# Mathematical-morphologyfunction [outputimgs] = morphology(filepath,rows,cols,bands,outputfile)
%MONORGRAPH 
%   contents
%Is image file exist?
if(exist(filepath,'file') == 0)
 error('image file unexist');
end
%Is hdr file exist?
hdrfile = strrep(filepath,'.dat','.hdr');
if(exist(hdrfile,'file') == 0)
 error('header file unexist');
end
outputimgs = zeros(rows,cols,bands);
imgs = multibandread(filepath,[rows,cols,bands],'single',0,'bsq','ieee-le');
%open first then close
for i=1:bands
    se = strel('square',3);%using square shape(3*3)
    img = imopen(imgs(:,:,i),se);%open first
    outputimgs(:,:,i) = imclose(img,se);%then close
end
%output image file
multibandwrite(outputimgs,outputfile,'bsq','precision','single','offset',0,'machfmt','ieee-le');
outputhdtfile = strrep(outputfile,'.dat','.hdr');
%generate hdr file in response to output file
copyfile(hdrfile, outputhdtfile);
end

